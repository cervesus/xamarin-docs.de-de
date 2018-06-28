---
title: Installieren der Xamarin-Vorschauversion unter Windows
description: In diesem Artikel wird beschrieben, wie Sie eine Vorschauversion von Xamarin in Visual Studio 2017 installieren, indem Sie den Kanal für die Veröffentlichung von Vorschauversionen verwenden.
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: asb3993
ms.author: amburns
ms.date: 03/20/2018
ms.openlocfilehash: d920dd688a7911ccf4002d67914c977da56f89e1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780684"
---
# <a name="installing-xamarin-preview-on-windows"></a>Installieren der Xamarin-Vorschauversion unter Windows

Wie in den vorherigen Versionen werden die Alpha- und die Betaversion sowie stabile Kanäle nicht von Visual Studio 2017 unterstützt. Es gibt stattdessen nur zwei Möglichkeiten:

- **Release**: entspricht dem _stabilen_ Kanal in Visual Studio für Mac
- **Vorschauversion**: entspricht den _Alpha_-, und _Beta_-Kanälen in Visual Studio für Mac

> [!TIP] 
> Um Vorabfeatures auszuprobieren, können Sie die Option [Download the Visual Studio 2017 Preview installer](https://www.visualstudio.com/vs/preview/) (Installer für Visual Studio 2017-Vorschauversion herunterladen) wählen. Damit können Sie **Vorschauversionen** von Visual Studio parallel zur stabilen (Vorschau-)Version installieren. Weitere Informationen zu Neuerungen in Visual Studio 2017 finden Sie in den [Versionsanmerkungen](/visualstudio/releasenotes/vs2017-preview-relnotes).

Die Vorschauversion von Visual Studio enthält möglicherweise die zugehörigen Vorschauversionen von Xamarin-Funktionen. Zu diesen gehören:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Workbooks/Inspector
- Xamarin Remote iOS Simulator

Der folgende Screenshot des **Vorschauversionsinstallers** zeigt sowohl Optionen für die Vorschauversion als auch für das Release (beachten Sie die grauen Versionsnummern: Version 15.0 ist das Release und Version 15.1 ist die Vorschauversion):

![Installer mit Optionen für die Vorschauversion](windows-images/vs2017-installer.jpg)

Wie im Folgenden dargestellt, kann während der Installation ein **Installationsspitzname** für die parallele Installation festgelegt werden (sodass diese im Startmenü voneinander unterschieden werden können):

[![Spitzname vor der Installation bearbeiten](windows-images/vs2017-nickname-sml.png "edit nickname before installing")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2017-preview"></a>Deinstallieren von Visual Studio 2017 Preview

Der **Visual Studio-Installer** sollte auch zur Deinstallation von Vorschauversionen von Visual Studio 2017 verwendet werden. Weitere Informationen finden Sie im [Leitfaden zum Deinstallieren von Xamarin](uninstalling-xamarin.md#uninstallvs2017).
