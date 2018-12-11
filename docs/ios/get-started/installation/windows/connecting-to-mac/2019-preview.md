---
title: Verbinden von Vorschauversionen von Visual Studio 2019 unter Windows und macOS
description: Dieses Handbuch enthält Anweisungen zur Verwendung der Vorschauversion von Visual Studio 2019 unter Windows zum Erstellen von iOS-Anwendungen, und die Vorschau von Visual Studio 2019 für Mac unter macOS wird zum Hosten Ihrer Builds verwendet.
ms.prod: xamarin
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/04/2018
ms.openlocfilehash: 1eeb79737bd712da64ce34a6b4fe83ce2823e3eb
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899363"
---
# <a name="visual-studio-2019-preview-pairing"></a>Kopplung der Vorschauversion von Visual Studio 2019

![Vorschau](~/media/shared/preview.png)

Bei der Vorschauversion von [Visual Studio für Mac 2019](https://docs.microsoft.com/visualstudio/mac/install-preview) wird die parallele Installation von Visual Studio auf MacOS erstmals vollständig unterstützt. Dies schafft eine neue Situation für Windows-Entwickler, die iOS-Anwendungen erstellen: Wenn ihr Mac-Buildhost sowohl Visual Studio 2017 für Mac (Version 7.x) als auch die Vorschauversion von Visual Studio 2019 für Mac (Version 8.0) enthält, welche Version agiert dann als Buildhost für ihre Windows Installation?

_Standardmäßig_ wird das stabile Release &ndash;Visual Studio 2017 für Mac (Version 7.7) &ndash; von Visual Studio unter Windows verwendet, unabhängig davon, ob Sie Visual Studio 2017 oder die Vorschauversion von Visual Studio 2019 unter Windows verwenden.

So testen Sie die Vorschauversionen von Visual Studio 2019 sowohl unter Windows als auch unter macOS ordnungsgemäß:

1. Installieren und verwenden Sie die Vorschauversion von Visual Studio 2019 (Version 16.0) auf Ihrem Windows-Computer.
2. Installieren Sie die Vorschauversionen von Visual Studio 2019 für Mac (Version 8.0) auf Ihrem Mac.
3. In **Visual Studio 2019 für Mac**:

    a. Wählen Sie das Menüelement **Visual Studio > Nach Updates suchen** aus.

    b. Wählen Sie im Fenster **Visual Studio-Update** die Option **Xamarin-Vorschauversion** aus dem Updatekanal. Die folgende Warnung wird angezeigt:

    > [!WARNING]
    > Testen Sie die neuesten Visual Studio 2019 für Mac Vorschau-Builds mit den neuesten Mono Runtime und Xamarin SDKs. **Die Installation von Builds aus diesem Kanal führt zur Unterbrechung des Visual Studio 2017 für Mac-Release.** Verwenden Sie diesen Kanal nur, wenn Sie Xamarin-Anwendungen auf einem Windows-Betriebssystem mit den Visual Studio 2019-Vorschauversionen erstellen.

    c. Drücken Sie die Schaltfläche **Kanal wechseln**, um den Buildhost der Vorschauversion zu aktivieren.

4. Befolgen Sie die Anweisungen unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](index.md) und [Tipps zur Problembehandlung](troubleshooting.md) (falls erforderlich).