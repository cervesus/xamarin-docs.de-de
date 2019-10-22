---
title: Quelllink mit xamarin. Forms
description: In diesem Artikel wird erläutert, wie Sie den quelllink zum Debuggen in xamarin. Forms verwenden.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetId: 1E13FCD9-5607-46E8-80E4-87A58B389BEB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/26/2019
ms.openlocfilehash: 7a3fe70c8ac29f9e84b5d071a0ba1ef73afbbe11
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697806"
---
# <a name="source-link-with-xamarinforms"></a>Quelllink mit xamarin. Forms

Xamarin. Forms-nuget-Pakete enthalten Zuordnungen von Quell Verknüpfungen. Mit der Quell Verknüpfung werden kompilierte Bibliotheken, die in einem nuget-Paket enthalten sind, einem Quellcoderepository zugeordnet. Visual Studio lädt Quell Code Dateien während des Debuggens herunter und ermöglicht es Entwicklern, Code schrittweise zu durchlaufen und so das Debuggen von Paketen zu ermöglichen, ohne

Weitere Informationen zum Verwenden des Quell Links finden Sie in der [Dokumentation zum quelllink](/dotnet/standard/library-guidance/sourcelink).

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019 unterstützt den quelllink für den **.NET-Debugger** , unterstützt derzeit jedoch keinen quelllink für den **Mono-Debugger**. Daher können Sie den quelllink zum Debuggen von UWP-Apps verwenden, jedoch nicht für die Android-oder IOS-app. Wenn Sie UWP-apps Debuggen, müssen Sie sicherstellen, dass die PDB-Dateien für Bibliotheken, die Sie debuggen möchten, in den Ordner " **AppX** " im Verzeichnis " **bin** " kopiert werden, in

## <a name="enable-source-link"></a>Quelllink aktivieren

Die Verwendung des Quell Links erfordert das Aktivieren des Debuggens für externen Code; andernfalls führt der Debugger den Code, der nicht in der aktuellen Projekt Mappe enthalten ist, aus. In Visual Studio 2019 finden Sie diese im Menü " **Optionen** " im Abschnitt " **Debuggen** ":

[![Enable Quell Verknüpfung in Visual Studio 2019](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

Stellen Sie sicher, dass " **nur eigenen Code aktivieren** " deaktiviert ist und dass **Unterstützung für quelllink aktivieren** aktiviert ist.

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>Quelllink aktivieren

Die Verwendung des Quell Links erfordert das Aktivieren des Debuggens für externen Code; andernfalls führt der Debugger den Code, der nicht in der aktuellen Projekt Mappe enthalten ist, aus. Diese Option finden Sie im Fenster " **Einstellungen** " im Abschnitt " **Debugger** ":

[![Enable Quell Verknüpfung in Visual Studio für Mac](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

Stellen Sie sicher, dass **externer Code in Einzelschritt** aktiviert ist.

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Debuggen von xamarin. Forms mithilfe des Quell Links

Wenn das Debuggen externer Pakete aktiviert ist, verwendet Visual Studio die Quell Link Zuordnungen, die im nuget-Paket enthalten sind, um den externen Quellcode herunterzuladen und schrittweise zu durchlaufen. Dies kann getestet werden, indem ein Haltepunkt beim Abrufen einer Methode festgelegt wird, die von xamarin. Forms bereitgestellt wird:

[![Breakpoint für die xamarin. Forms-Methode festgelegt.](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Abhängig von den Einstellungen, die Sie in den **Debuggeroptionen** angegeben haben, werden Sie von Visual Studio gewarnt, dass Quelldateien heruntergeladen werden:

[Warnung bei externem Code in ![Visual Studio](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Wenn Sie zulassen, dass Visual Studio die Dateien herunterlädt, führt der Debugger einen Einzelschritt in den externen Code aus.

::: zone pivot="windows"

## <a name="source-link-caching"></a>Zwischenspeichern von Quell Links

Quell Link verwendet Caching für die Leistung. Das cachingverzeichnis für den quelllink wird im Menü " **Optionen** " unter " **Debuggen** " im Abschnitt " **Symbole** " definiert:

[Zwischenspeichern von ![Visual Studio-Quell Links](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

Mit diesem Menü können Sie das cachingverzeichnis für alle Debugsymbole angeben und den Cache löschen, wenn Sie Probleme mit zwischengespeicherten Symbolen haben.

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>Zwischenspeichern von Quell Links

Quell Link verwendet Caching für die Leistung. Das Caching-Verzeichnis für den quelllink unter MacOS ist `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols`. Dieser Ordner enthält Unterordner, in denen das Repository zum Herunterladen von Quelldateien gespeichert wird. Wenn sich das Unterstützungs Repository für ein nuget-Paket geändert hat, müssen Sie diese Ordner möglicherweise manuell löschen, um den Cache zu aktualisieren.

::: zone-end

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zum quelllink](/dotnet/standard/library-guidance/sourcelink)
- [Quelllink auf GitHub](https://github.com/dotnet/sourcelink)
