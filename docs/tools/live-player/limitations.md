---
title: Einschränkungen
description: Einige Einschränkungen von der Xamarin-Live-Player
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: 71d37b79c962123bb5adf6f999e713f68086166c
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="limitations"></a>Einschränkungen

![Vorschaufunktion](~/media/shared/preview.png)

## <a name="device-requirements"></a>Geräteanforderungen
Die Live-Player Xamarin-app unterstützt die folgenden Geräte:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 oder höher.
- ARM-v7a, ARM-v8a, ARM64-v8a, x 86- oder x86_64-Prozessor.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 oder höher.
- ARM64-Prozessor.

-----

## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen auf die Dinge, die Xamarin-Live-Player ausgeführt werden kann, einschließlich der folgenden Elemente:

### <a name="xamarinforms"></a>Xamarin.Forms
- Benutzerdefinierte Renderer werden nicht unterstützt.
- Effekte werden nicht unterstützt.
- Benutzerdefinierte Steuerelemente mit benutzerdefinierten bindbare Eigenschaften werden nicht unterstützt.
- Eingebettete Ressourcen werden nicht unterstützt (d. h. Einbetten von Bildern oder anderen Ressourcen in einer PCL).
- MVVM-Frameworks von Drittanbietern werden (also nicht unterstützt. Prism, Mvvm Cross, Mvvm Light, usw.).
- Asset-Kataloge unter iOS werden nicht unterstützt.

### <a name="other-project-types"></a>Andere Projekttypen
- Live-Player ist nicht vorgesehen, für systemeigenes Android oder iOS-Projekte (die Android-XML-Index oder Storyboards für die Benutzeroberfläche zu verwenden).

### <a name="misc"></a>Sonst.
- Eingeschränkte Unterstützung für die Reflektion (derzeit einige beliebte NuGets, z. B. SQLite und Json.NET betrifft). Andere NuGets möglicherweise immer noch unterstützt werden.
- Einige Systemklassen nicht überschrieben werden kann (z. B. eine Unterklasse kann nicht implementiert werden).
- Einige Plattformfunktionen, die Bereitstellung erforderlich, können nicht in der Live-Player Xamarin-app (jedoch er bei allgemeinen Vorgängen wie Foto-Katalog-Zugriff konfiguriert wurde) funktionieren.
- Benutzerdefinierte Ziele und Buildschritte werden ignoriert. Z. B. Tools wie Fody, Retit, AutoFac und AutoMapper kann nicht integriert werden.
- F#-Projekten werden nicht von Android unterstützt und eingeschränkte Unterstützung für iOS
- Erweiterte Szenarien mit benutzerdefinierten generischen Klassen und Schnittstellen wird möglicherweise nicht unterstützt.

Melden Sie alle weiteren Probleme auf [Bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
