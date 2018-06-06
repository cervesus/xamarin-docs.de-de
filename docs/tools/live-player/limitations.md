---
title: Einschränkungen von Xamarin Live Player
description: Dieses Dokument beschreibt die Einschränkungen von der Xamarin-Live-Player. Es erläutert die geräteanforderungen, bietet es funktioniert mit Projekttypen und anderen verschiedenen Themen.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793674"
---
# <a name="limitations-of-xamarin-live-player"></a>Einschränkungen von Xamarin Live Player

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
- Benutzerdefinierte Ziele und Buildschritte werden ignoriert. Tools wie Fody, rüsten AutoFac und AutoMapper können z. B. können nicht integriert werden.
- F#-Projekten werden nicht von Android unterstützt und eingeschränkte Unterstützung für iOS
- Erweiterte Szenarien mit benutzerdefinierten generischen Klassen und Schnittstellen wird möglicherweise nicht unterstützt.

Melden Sie alle weiteren Probleme auf [Bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
