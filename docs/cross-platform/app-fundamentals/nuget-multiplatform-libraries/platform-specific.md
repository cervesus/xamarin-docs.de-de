---
title: Erstellen neue Projekte der plattformspezifischen Bibliothek für NuGet
description: Dieses Dokument beschreibt, wie Sie ein einzelnes NuGet-Paket zu erstellen, das enthält plattformspezifischen Code für mehrere Plattformen.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4be010448d963462ccf06c263ddfad7ba1d9feae
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832036"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Erstellen neue Projekte der plattformspezifischen Bibliothek für NuGet

Für mehrere Plattformen Library-Projekten, die auf bestimmte Plattformen wie iOS und Android ausgerichtet funktionieren am besten mit freigegebenen Projekten.

NuGet kann sowohl iOS und Android-spezifischen Code, als auch .NET Code sowohl enthalten.

Mehrere Assemblys erstellt und in einem einzelnen NuGet-Paket integriert. NuGet-Standards wird sichergestellt, dass das Paket für alle unterstützten Projekttypen, z. B. Xamarin.iOS- und Android-Projekte hinzugefügt werden kann.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Schritte zum Erstellen von plattformübergreifenden Bibliothek NuGet

1. Wählen Sie **Datei > neue Projektmappe** (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projektmappe, und wählen Sie **hinzufügen > Neues Projekt**).

2. Wählen Sie **Multi-Plattform-Bibliothek** aus der **Multi-Plattform > Bibliothek** Abschnitt:

    [![](platform-specific-images/mulitplatform-library-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzigen Codebasis")](platform-specific-images/multiplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und **Beschreibung**, und wählen Sie **plattformspezifischen**:

    [![](platform-specific-images/specific-configure-sml.png "Konfigurieren der plattformspezifischen Bibliothek für iOS und Android")](platform-specific-images/specific-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. Die folgenden Projekte werden der Projektmappe hinzugefügt:

    - **Android-Projekt** – Android-spezifischem Code kann optional an diesem Projekt hinzugefügt werden.
    - **iOS-Projekts** – iOS-spezifischem Code kann optional an diesem Projekt hinzugefügt werden.
    - **NuGet-Projekt** – kein Code zu diesem Projekt hinzugefügt. Es verweist auf die anderen Projekte, und die Metadatenkonfiguration für die Ausgabe der NuGet-Paket enthält.
    - **Freigegebenes Projekt** – gemeinsamer Code einschließlich der plattformspezifischen Code in diesem Projekt hinzugefügt werden sollen `#if` Compiler-Direktiven.

5. Mit der rechten Maustaste auf das NuGet-Projekt, und wählen Sie **Optionen**, und öffnen Sie dann die **NuGet-Pakets > Metadaten** aus, und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (als sowie alle optional die Metadaten):

    [![](platform-specific-images/specific-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](platform-specific-images/specific-metadata.png#lightbox)

6. Auch in der **Projektoptionen** geöffnete Fenster die **Verweisassemblys** aus, und wählen Sie die PCL-Profilen, die die freigegebene Bibliothek per "lockvogel" unterstützt:

    ![](platform-specific-images/specific-reference-assemblies.png "Öffnen Sie im Fenster \"Projektoptionen\" den Abschnitt der Referenz-Assemblys außerdem wählen Sie die PCL-Profilen, die die freigegebene Bibliothek über lockvogel unterstützt werden")

    > [!NOTE]
    > "Lockvogel" bedeutet, dass die PCL-Assemblys nur enthalten, werden die API, die von der Bibliothek (es kann keine der plattformspezifischen Code enthalten) verfügbar gemacht werden. Wenn NuGet ein Xamarin-Projekt hinzugefügt wird, gemeinsam genutzte Bibliotheken für die PCL kompiliert werden, aber die plattformspezifische Assemblys enthalten den Code, der tatsächlich von den IOS- oder Android-Projekt verwendet wird.

7. Mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Paket erstellen** (oder erstellen oder Bereitstellen der Lösung) und die **NUPKG-Datei** NuGet-Paket-Datei wird gespeichert der **/bin /** Ordner ( einer Debug- oder Releasekonfiguration, je nach Konfiguration).

    ![](platform-specific-images/create-nuget-package.png "NuGet-Paket-Datei wird im Ordner \"Bin\" einer Debug- oder Releasekonfiguration, je nach Konfiguration gespeichert werden")


## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch die ZIP-Dateien, daher ist es möglich, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines plattformspezifischen NuGet, die unterstützt iOS und Android und hatte zwei Verweisassemblys ausgewählt:

![](platform-specific-images/nuget-output.png "Im NuGet-Paket enthaltenen Dateien")


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
