---
title: "Erstellen eine neue Multiplattform-Bibliothek für NuGet"
ms.topic: article
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e95cf18c281732c85c2029e4ff35e8dd8be0f5e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Erstellen eine neue Multiplattform-Bibliothek für NuGet

Erstellen ein Klassenbibliotheksprojekt für mehrere Plattformen, die PCL verwendet oder .NET Standard bedeutet, dass die resultierende NuGet zu einem Projekt .NET kann, die das Zielprofil hinzugefügt werden, einschließlich ASP.NET-Projekte oder desktop-apps mit WinForms, WPF oder universelle Windows-Plattform unterstützt.

Die Bibliothek darf nur Code unterstützt das ausgewählte PCL "oder" Standard ".NET Profil als auch alle anderen NuGets, die hinzugefügt werden.
Dies eignet sich für die Geschäftslogik und Algorithmen, die vollständig in der Basisklassenbibliothek .NET ausgedrückt werden können.

Eine einzelne Assembly erstellt und in ein NuGet-Paket integriert.

Wenn Sie später Clientplattform-spezifische Funktionalität benötigen [plattformspezifischen Projekten hinzugefügt werden können](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Schritte zum Erstellen einer Bibliothek für mehrere Plattformen NuGet

1. Wählen Sie **Datei > neue Projektmappe** (oder klicken Sie mit der rechten Maustaste auf einer vorhandenen Projektmappe, und wählen Sie **hinzufügen > Neues Projekt**).

2. Wählen Sie **Bibliothek für mehrere Plattformen** aus der **mehrere Plattformen > Bibliothek** Abschnitt:

  [ ![](single-codebase-images/mulitplatform-library-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzelnen Codebasis")](single-codebase-images/mulitplatform-library.png)

3. Geben Sie einen **Namen** und **Beschreibung**, und wählen Sie **für alle Plattformen einzelne**:

  [ ![](single-codebase-images/single-configure-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzelnen Codebasis")](single-codebase-images/single-configure.png)

4. Durchlaufen Sie den Assistenten. Eine einzelne Library-Projekt wird in der Projektmappe erstellt.

5. Mit der rechten Maustaste auf das neue Bibliotheksprojekt, und wählen Sie dann **Optionen**. Die **erstellen > Allgemein** Abschnitt ermöglicht den **Zielframework** festgelegt werden: Wählen Sie ein .NET Portable PCL-Profil oder eine .NET Standardversion:

  [ ![](single-codebase-images/single-choose-type-sml.png "Wählen Sie für Bibliothekstyp PCL "oder" Standard ".NET")](single-codebase-images/single-choose-type.png)

6. Auch in der **Projektoptionen** geöffnete Fenster die **NuGet-Paket > Metadaten** Abschnitt, und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten):

  [ ![](single-codebase-images/single-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](single-codebase-images/single-metadata.png)

7. Mit der rechten Maustaste auf das Steuerelementbibliothek-Projekt, und wählen Sie **NuGet-Pakete erstellen** (oder erstellen oder Bereitstellen der Projektmappe) und die **.nupkg** NuGet-Paket-Datei wird gespeichert, der **/bin/** Ordner (einer Debug- oder Releasekonfiguration, je nach Konfiguration):

  ![](single-codebase-images/create-nuget-package.png "Die NuGet-Paket-Datei wird im Ordner "Bin" einer Debug- oder Releasekonfiguration, je nach Konfiguration gespeichert werden")


## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch ZIP-Dateien, daher ist es möglich, die interne Struktur der generierten Paket zu überprüfen.

Diese bildschirmabbildung zeigt den Inhalt von einem PCL-basierte NuGet – nur eine einzelne PCL-Assembly enthalten ist:

![](single-codebase-images/nuget-output.png "Dateien in das NuGet-Paket")

<a name="add-platforms" />

# <a name="adding-platform-specific-code"></a>Hinzufügen von plattformspezifischen Code

PCL-basierten und .NET Standard-basierte Projekte können keine plattformspezifischen Verweise (z. B. iOS oder Android-Funktionalität) enthalten.

Wenn eine vorhandene PCL-Projekt oder ein .NET Standard Projekt werden, um plattformspezifischen Code enthalten erweitert muss, dies kann erfolgen, indem mit der rechten Maustaste auf das Projekt klicken und auswählen **hinzufügen > Plattform Implementierung hinzufügen...** :

[ ![](single-codebase-images/add-later-sml.png "Plattform Implementierung Menü "hinzufügen"")](single-codebase-images/add-later.png)

Eine oder mehrere plattformprojekten können der Projektmappe hinzugefügt werden, und die vorhandene PCL "oder" Standard ".NET Bibliothek optional zu einem freigegebenen Projekt konvertiert werden kann:

[ ![](single-codebase-images/add-later-platforms-sml.png "Optionen für Plattformen wie iOS, Android und freigegebenes Projekt hinzufügen")](single-codebase-images/add-later-platforms-sml.png)

Nach der Konvertierung in ein freigegebenes Projekt, besuchen Sie die **Projektoptionen > NuGet-Paket > Verweisassemblys**
[Abschnitt](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) und stellen Sie sicher, dass alle erforderlichen Profile ausgewählt werden (so, dass die NuGet weiterhin mit Projekten kompatibel sein, die im zuvor verwendet wurde).


## <a name="related-links"></a>Verwandte Links

- [Metadaten-Handbuch](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
