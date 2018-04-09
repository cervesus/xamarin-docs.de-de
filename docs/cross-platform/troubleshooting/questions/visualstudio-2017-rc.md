---
redirect_url: /xamarin/cross-platform/troubleshooting/questions/
title: Kann ich Visual Studio 2017 Release Candidate mit Xamarin verwenden?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cfad562bfbfbc3985efa6252aa8eb9b6559fcc41
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Kann ich Visual Studio 2017 Release Candidate mit Xamarin verwenden?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Kann ich Visual Studio 2017 Release Candidate mit Xamarin verwenden?

Ja. Sie sind über Visual Studio 2017 Release Candidate Xamarin nutzen können. Beachten Sie jedoch, dass derzeit früheren Versionen von Xamarin in Visual Studio 2015/2013 installiert Installieren von Xamarin für Visual Studio 2017 RC deinstalliert wird. Xamarin für Visual Studio-2017 kann nicht zur gleichen Zeit wie Xamarin für Visual Studio 2015/2013 als Ergebnis der Visual Studio 2017 Verschieben von MSI-Paket in Richtung der Auslastung des Systems Installer für Visual Studio installiert werden.

Während das Team derzeit in Möglichkeiten sucht, dieses Verhalten zu umgehen, wir empfehlen, ihre Entwicklungsumgebung, die je nach ihren Anforderungen auszuwählen. 

> [!IMPORTANT]
> Wenn Sie Xamarin.iOS Projekte erstellen, stellen Sie sicher, dass Visual Studio für Mac in Ihrem paarweise zugeordneten Mac-System auf die gleiche Version von Xamarin.iOS Kanal ist.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Wie installiere ich Xamarin in Visual Studio 2017 Release Candidate?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Während der 2017 RC Installer für Visual Studio installieren

* Wählen Sie die **Xamarin** -Komponente als Teil der neuen **Installer für Visual Studio**

  [![](visualstudio-2017-rc-images/install1-sml.png "Visual Studio 2017 RC Installer-Bildschirm")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

Hiermit wird der Visual Studio-Erweiterung für die Entwicklung von Xamarin.iOS und Xamarin.Android installiert.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Installieren oder eine erneute Installation von Xamarin in einer vorhandenen Installation von Visual Studio 2017 RC

#### <a name="using-the-visual-studio-installer"></a>Verwenden die Visual Studio-Installer:

1. Suchen Sie nach der Installer für Visual Studio-Anwendung

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Suchergebnisse für Visual Studio Installer-Anwendung")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. Wählen Sie: ein. **Mobile Entwicklung mit .NET (Vorschau)** auf der Registerkarte Arbeitslasten oder

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "Registerkarte "Visual Studio Installer-Arbeitslasten"") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** in der **Einzelkomponenten** Registerkarte

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "Registerkarte "Visual Studio Installer-Komponenten"")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Verwenden den Installer für Visual Studio innerhalb von Visual Studio:
1. Navigieren Sie zu Visual Studio 2017-Startseite
2. Klicken Sie auf **mehr Projektvorlagen** unter der **neues Projekt** Abschnitt

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Visual Studio-Startseite")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. Klicken Sie auf `Open Visual Studio Installer` im linken Bereich

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "Bildschirm "Neues Projekt"")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. Wählen Sie Folgendes aus:
    
    a. **Mobile Entwicklung mit .NET (Vorschau)** auf der Registerkarte Arbeitslasten oder

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "Registerkarte "Visual Studio Installer-Arbeitslasten"") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** in der **Einzelkomponenten** Registerkarte

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "Registerkarte "Visual Studio Installer-Komponenten"")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
