---
title: 'title: "Installieren der Xamarin-Vorschauversion unter Windows" description: "In diesem Artikel wird beschrieben, wie Sie eine Vorschauversion von Xamarin in Visual Studio 2019 installieren, indem Sie den Kanal für die Veröffentlichung von Vorschauversionen verwenden."'
description: 'ms.prod: xamarin ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA author: conceptdev ms.author: crdun ms.date: 03/20/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ccd5a610ad41c0160a6778a63a367376bd200b3
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84134016"
---
# <a name="installing-xamarin-preview-on-windows"></a>Installieren der Xamarin-Vorschauversion unter Windows

Wie in den vorherigen Versionen werden die Alpha- und die Betaversion sowie stabile Kanäle nicht von Visual Studio 2019 und Visual Studio 2017 unterstützt. Es gibt stattdessen nur zwei Möglichkeiten:

- **Release**: entspricht dem _stabilen_ Kanal in Visual Studio für Mac
- **Vorschauversion**: entspricht den _Alpha_-, und _Beta_-Kanälen in Visual Studio für Mac

> [!TIP]
> Um Vorabfeatures auszuprobieren, können Sie die Option [Download the Visual Studio Preview installer](https://visualstudio.microsoft.com/vs/preview/) (Installer für Visual Studio-Vorschauversion herunterladen) auswählen. Damit können Sie **Vorschauversionen** von Visual Studio parallel zur stabilen Vorschauversion installieren. Weitere Informationen zu Neuerungen in Visual Studio 2019 finden Sie in den [Versionshinweisen](https://docs.microsoft.com/visualstudio/releases/2019/release-notes).

Die Vorschauversion von Visual Studio enthält möglicherweise die zugehörigen Vorschauversionen von Xamarin-Funktionen. Zu diesen gehören:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Inspector
- Xamarin Remote iOS Simulator

Der folgende Screenshot des **Vorschauversionsinstallers** zeigt sowohl Optionen für die Vorschauversion als auch für das Release (beachten Sie die grauen Versionsnummern: Version 15.0 ist das Release und Version 15.1 ist die Vorschauversion):

![Installer mit Optionen für die Vorschauversion](windows-images/vs2017-installer.jpg)

Wie im Folgenden dargestellt, kann während der Installation ein **Installationsspitzname** für die parallele Installation festgelegt werden (sodass diese im Startmenü voneinander unterschieden werden können):

[![Bearbeitung des Spitznamens vor der Installation](windows-images/vs2017-nickname-sml.png "Bearbeitung des Spitznamens vor der Installation")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2019-preview"></a>Deinstallieren von Visual Studio 2019 Preview

Der **Visual Studio-Installer** sollte auch zur Deinstallation von Vorschauversionen von Visual Studio 2019 verwendet werden. Weitere Informationen finden Sie im [Leitfaden zum Deinstallieren von Xamarin](uninstalling-xamarin.md#uninstallvs2017).
