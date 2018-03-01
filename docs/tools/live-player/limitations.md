---
title: "Einschränkungen"
description: "Einige Einschränkungen von der Xamarin-Live-Player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Einschränkungen

![Vorschaufunktion](~/media/shared/preview.png)

## <a name="device-requirements"></a>Geräteanforderungen
Die Live-Player Xamarin-app unterstützt die folgenden Geräte:

### <a name="ios"></a>iOS
- iOS 9.0 oder höher.
- ARM64-Prozessor.
- Überprüfen Sie die [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) eine Liste der unterstützten Geräte.

### <a name="android"></a>Android
- Android 4.2 oder höher.
- ARM-v7a, ARM-v8a, ARM64-v8a, x 86- oder x86_64-Prozessor.


## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen auf die Dinge, die Xamarin-Live-Player ausgeführt werden kann, einschließlich der folgenden Elemente:

### <a name="android"></a>Android
- Android-Benutzer-Schnittstellen, die mit AXML Dateien konzipiert werden derzeit nicht unterstützt.

### <a name="ios"></a>iOS
- Einige iOS-Storyboard-Funktionen werden nicht unterstützt.
- iOS XIB Dateien werden nicht unterstützt.

### <a name="xamarinforms"></a>Xamarin.Forms
- Benutzerdefinierte Renderer werden nicht unterstützt.
- Effekte werden nicht unterstützt.
- Benutzerdefinierte Steuerelemente mit benutzerdefinierten bindbare Eigenschaften werden nicht unterstützt.
- Eingebettete Ressourcen werden nicht unterstützt (d. h. Einbetten von Bildern oder anderen Ressourcen in einer PCL).

### <a name="misc"></a>Sonstiges
- Eingeschränkte Unterstützung für die Reflektion (derzeit einige beliebte NuGets, z. B. SQLite und Json.NET betrifft). Andere NuGets werden weiterhin unterstützt.
- Einige Systemklassen nicht überschrieben werden kann (z. B. eine Unterklasse kann nicht implementiert werden).
- Einige Plattformfunktionen, die Bereitstellung erforderlich, können nicht in der Live-Player Xamarin-app (jedoch er bei allgemeinen Vorgängen wie Foto-Katalog-Zugriff konfiguriert wurde) funktionieren.
- Benutzerdefinierte Ziele und Buildschritte werden ignoriert. Dieser Tools z. B. ein, wie z. B. Fody integriert werden kann.

> [!WARNING]
> **Hinweis:** Xamarin Live Player funktioniert nicht mit Xamarin Studio

Melden Sie alle weiteren Probleme auf [Bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
