---
title: Erstellen eine neue plattformübergreifende Bibliothek für NuGet
description: Dieses Dokument beschreibt, wie Sie eine plattformübergreifende Bibliothek für die Verwendung mit NuGet zu erstellen. Dieses Verfahren eignet sich für die Geschäftslogik und Algorithmen, die vollständig in der Klassenbibliothek von .NET Base ausgedrückt werden können und daher auf allen Zielplattformen ohne plattformspezifische Code ausgeführt werden.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6d695df9c59a5f95441092d6d7b44d5feda941bd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61267758"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Erstellen eine neue plattformübergreifende Bibliothek für NuGet

Erstellen ein Multi-Plattform-Bibliotheksprojekt, das PCL verwendet oder .NET Standard bedeutet, dass die resultierende NuGet zu jedem .NET-Projekt kann, die das Zielprofil hinzugefügt werden, z. B. ASP.NET-Projekte oder desktop-apps mithilfe von WinForms, WPF oder UWP unterstützt.

Enthält die Bibliothek kann nur Code unterstützt durch das ausgewählte Profil für PCL- oder .NET Standard als auch alle anderen NuGet-Pakete, die hinzugefügt werden.
Dies ist geeignet, Geschäftslogik und Algorithmen, die vollständig in der Basisklassenbibliothek von .NET ausgedrückt werden können.

Eine einzelne Assembly erstellt und in ein NuGet-Paket integriert.

Wenn Sie später plattformspezifische Funktionalität benötigen [plattformspezifischen Projekten hinzugefügt werden können](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Schritte zum Erstellen eines NuGet-Bibliothek für mehrere Plattformen

1. Wählen Sie **Datei > neue Projektmappe** (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projektmappe, und wählen Sie **hinzufügen > Neues Projekt**).

2. Wählen Sie **Multi-Plattform-Bibliothek** aus der **Multi-Plattform > Bibliothek** Abschnitt:

  [![](single-codebase-images/mulitplatform-library-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzigen Codebasis")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und **Beschreibung**, und wählen Sie **einmal für alle Plattformen**:

  [![](single-codebase-images/single-configure-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzigen Codebasis")](single-codebase-images/single-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. Eine einzelne Library-Projekt wird in der Projektmappe erstellt.

5. Mit der rechten Maustaste auf das neue Bibliotheksprojekt, und wählen Sie dann **Optionen**. Die **erstellen > Allgemein** Abschnitt können die **Zielframework** festzulegende – wählen Sie ein .NET Portable-PCL-Profil oder .NET Standard-Version:

  [![](single-codebase-images/single-choose-type-sml.png "Wählen Sie für den Typ \"Klassenbibliothek\" PCL- oder .NET Standard")](single-codebase-images/single-choose-type.png#lightbox)

6. Auch in der **Projektoptionen** geöffnete Fenster die **NuGet-Paket > Metadaten** aus, und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten):

  [![](single-codebase-images/single-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](single-codebase-images/single-metadata.png#lightbox)

7. Mit der rechten Maustaste auf das Bibliotheksprojekt, und wählen Sie **NuGet-Paket erstellen** (oder erstellen oder Bereitstellen der Lösung) und die **NUPKG-Datei** NuGet-Paket-Datei wird gespeichert der **/bin /** Ordner (Debug oder Release, je nach Konfiguration):

  ![](single-codebase-images/create-nuget-package.png "Die NuGet-Paket-Datei wird im Ordner \"Bin\" einer Debug- oder Releasekonfiguration, je nach Konfiguration gespeichert werden")


## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch die ZIP-Dateien, daher ist es möglich, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten NuGet – nur eine einzelne PCL-Assembly enthalten ist:

![](single-codebase-images/nuget-output.png "Im NuGet-Paket enthaltenen Dateien")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Hinzufügen von plattformspezifischem Code

PCL-basierten Projekten und .NET Standard-basierte Projekte dürfen keine plattformspezifischen Referenzen (z. B. iOS oder Android-Funktionalität) enthalten.

Wenn ein vorhandenes PCL-Projekt oder die .NET Standard-Projekt werden, um plattformspezifischen Code einschließen erweitert muss, dies kann erfolgen, indem Sie mit der rechten Maustaste auf das Projekt, und wählen **hinzufügen > Hinzufügen der Plattformimplementierung...** :

[![](single-codebase-images/add-later-sml.png "Plattform-Implementierung-Menü \"hinzufügen\"")](single-codebase-images/add-later.png#lightbox)

Mindestens eine Plattform-Projekten können die Lösung hinzugefügt werden, und die vorhandene PCL- oder .NET Standard-Bibliothek in ein freigegebenes Projekt optional konvertiert werden kann:

[![](single-codebase-images/add-later-platforms-sml.png "Hinzufügen von Optionen für die Plattformen wie iOS, Android und freigegebenes Projekt")](single-codebase-images/add-later-platforms-sml.png#lightbox)

Besuchen Sie nach dem Konvertieren in ein freigegebenes Projekt, die **Projektoptionen > NuGet-Paket > Verweisassemblys**
[Abschnitt](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) und stellen Sie sicher, dass alle erforderlichen Profilen ausgewählt werden (so, dass die NuGet ist weiterhin mit Projekten kompatibel, die sie zuvor in verwendet wurde).


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
